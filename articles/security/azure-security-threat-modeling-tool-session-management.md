---
title: "Relace správy – nástroj Microsoft Threat modelování – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb, které jsou zveřejněné v nástroji pro modelování hrozeb"
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
ms.openlocfilehash: 56471d8ef68eacacb3ecebad5056d7e7a9f3ca40
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="security-frame-session-management--articles"></a><span data-ttu-id="c1b38-103">Rámce zabezpečení: Správa relací | Články</span><span class="sxs-lookup"><span data-stu-id="c1b38-103">Security Frame: Session Management | Articles</span></span> 
| <span data-ttu-id="c1b38-104">Produktům a službám</span><span class="sxs-lookup"><span data-stu-id="c1b38-104">Product/Service</span></span> | <span data-ttu-id="c1b38-105">Článek</span><span class="sxs-lookup"><span data-stu-id="c1b38-105">Article</span></span> |
| --------------- | ------- |
| <span data-ttu-id="c1b38-106">**Azure AD**</span><span class="sxs-lookup"><span data-stu-id="c1b38-106">**Azure AD**</span></span>    | <ul><li>[<span data-ttu-id="c1b38-107">Implementace správné odhlášení pomocí ADAL metod při používání služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1b38-107">Implement proper logout using ADAL methods when using Azure AD</span></span>](#logout-adal)</li></ul> |
| <span data-ttu-id="c1b38-108">Zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="c1b38-108">IoT Device</span></span> | <ul><li>[<span data-ttu-id="c1b38-109">Použití omezené životnost generovaných tokenů SaS</span><span class="sxs-lookup"><span data-stu-id="c1b38-109">Use finite lifetimes for generated SaS tokens</span></span>](#finite-tokens)</li></ul> |
| <span data-ttu-id="c1b38-110">**Azure Documentdb**</span><span class="sxs-lookup"><span data-stu-id="c1b38-110">**Azure Document DB**</span></span> | <ul><li>[<span data-ttu-id="c1b38-111">Pomocí minimální životnosti tokenu pro generovaných tokenů prostředků</span><span class="sxs-lookup"><span data-stu-id="c1b38-111">Use minimum token lifetimes for generated Resource tokens</span></span>](#resource-tokens)</li></ul> |
| <span data-ttu-id="c1b38-112">**ADFS**</span><span class="sxs-lookup"><span data-stu-id="c1b38-112">**ADFS**</span></span> | <ul><li>[<span data-ttu-id="c1b38-113">Implementace správné odhlášení metodami WsFederation při použití služby AD FS</span><span class="sxs-lookup"><span data-stu-id="c1b38-113">Implement proper logout using WsFederation methods when using ADFS</span></span>](#wsfederation-logout)</li></ul> |
| <span data-ttu-id="c1b38-114">**Serveru identit**</span><span class="sxs-lookup"><span data-stu-id="c1b38-114">**Identity Server**</span></span> | <ul><li>[<span data-ttu-id="c1b38-115">Implementace správné odhlášení při použití serveru identit</span><span class="sxs-lookup"><span data-stu-id="c1b38-115">Implement proper logout when using Identity Server</span></span>](#proper-logout)</li></ul> |
| <span data-ttu-id="c1b38-116">**Webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="c1b38-116">**Web Application**</span></span> | <ul><li>[<span data-ttu-id="c1b38-117">Aplikace dostupné přes protokol HTTPS, musíte použít bezpečné soubory cookie</span><span class="sxs-lookup"><span data-stu-id="c1b38-117">Applications available over HTTPS must use secure cookies</span></span>](#https-secure-cookies)</li><li>[<span data-ttu-id="c1b38-118">Všechny aplikace http, na základě by měl určovat pouze pro definici souboru cookie http</span><span class="sxs-lookup"><span data-stu-id="c1b38-118">All http based application should specify http only for cookie definition</span></span>](#cookie-definition)</li><li>[<span data-ttu-id="c1b38-119">Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na webových stránkách ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c1b38-119">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>](#csrf-asp)</li><li>[<span data-ttu-id="c1b38-120">Nastavení relace dobu jeho existence nečinnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-120">Set up session for inactivity lifetime</span></span>](#inactivity-lifetime)</li><li>[<span data-ttu-id="c1b38-121">Implementace správné odhlášení z aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-121">Implement proper logout from the application</span></span>](#proper-app-logout)</li></ul> |
| <span data-ttu-id="c1b38-122">**Webové rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="c1b38-122">**Web API**</span></span> | <ul><li>[<span data-ttu-id="c1b38-123">Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c1b38-123">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>](#csrf-api)</li></ul> |

## <span data-ttu-id="c1b38-124"><a id="logout-adal"></a>Implementace správné odhlášení pomocí ADAL metod při používání služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1b38-124"><a id="logout-adal"></a>Implement proper logout using ADAL methods when using Azure AD</span></span>

| <span data-ttu-id="c1b38-125">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-125">Title</span></span>                   | <span data-ttu-id="c1b38-126">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-126">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-127">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-127">**Component**</span></span>               | <span data-ttu-id="c1b38-128">Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1b38-128">Azure AD</span></span> | 
| <span data-ttu-id="c1b38-129">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-129">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-130">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-130">Build</span></span> |  
| <span data-ttu-id="c1b38-131">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-131">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-132">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-132">Generic</span></span> |
| <span data-ttu-id="c1b38-133">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-133">**Attributes**</span></span>              | <span data-ttu-id="c1b38-134">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-134">N/A</span></span>  |
| <span data-ttu-id="c1b38-135">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-135">**References**</span></span>              | <span data-ttu-id="c1b38-136">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-136">N/A</span></span>  |
| <span data-ttu-id="c1b38-137">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-137">**Steps**</span></span> | <span data-ttu-id="c1b38-138">Pokud aplikace využívá přístupový token vydán Azure AD, by měly volat obslužné rutiny události odhlášení</span><span class="sxs-lookup"><span data-stu-id="c1b38-138">If the application relies on access token issued by Azure AD, the logout event handler should call</span></span> |

### <a name="example"></a><span data-ttu-id="c1b38-139">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-139">Example</span></span>
```C#
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a><span data-ttu-id="c1b38-140">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-140">Example</span></span>
<span data-ttu-id="c1b38-141">Relace uživatele ho měli zrušte také voláním metody Session.Abandon().</span><span class="sxs-lookup"><span data-stu-id="c1b38-141">It should also destroy user's session by calling Session.Abandon() method.</span></span> <span data-ttu-id="c1b38-142">Následující metoda ukazuje zabezpečené implementací odhlášení uživatele:</span><span class="sxs-lookup"><span data-stu-id="c1b38-142">Following method shows secure implementation of user logout:</span></span>
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

## <span data-ttu-id="c1b38-143"><a id="finite-tokens"></a>Použití omezené životnost generovaných tokenů SaS</span><span class="sxs-lookup"><span data-stu-id="c1b38-143"><a id="finite-tokens"></a>Use finite lifetimes for generated SaS tokens</span></span>

| <span data-ttu-id="c1b38-144">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-144">Title</span></span>                   | <span data-ttu-id="c1b38-145">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-145">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-146">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-146">**Component**</span></span>               | <span data-ttu-id="c1b38-147">Zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="c1b38-147">IoT Device</span></span> | 
| <span data-ttu-id="c1b38-148">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-148">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-149">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-149">Build</span></span> |  
| <span data-ttu-id="c1b38-150">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-150">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-151">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-151">Generic</span></span> |
| <span data-ttu-id="c1b38-152">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-152">**Attributes**</span></span>              | <span data-ttu-id="c1b38-153">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-153">N/A</span></span>  |
| <span data-ttu-id="c1b38-154">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-154">**References**</span></span>              | <span data-ttu-id="c1b38-155">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-155">N/A</span></span>  |
| <span data-ttu-id="c1b38-156">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-156">**Steps**</span></span> | <span data-ttu-id="c1b38-157">Vygeneruje ověřování pro službu Azure IoT Hub tokeny SaS by měl mít dobou vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="c1b38-157">SaS tokens generated for authenticating to Azure IoT Hub should have a finite expiry period.</span></span> <span data-ttu-id="c1b38-158">Zachovat životnosti tokenu SaS s minimální omezit množství času, který může být přehrány v případě, že dojde k ohrožení tokenů.</span><span class="sxs-lookup"><span data-stu-id="c1b38-158">Keep the SaS token lifetimes to a minimum to limit the amount of time they can be replayed in case the tokens are compromised.</span></span>|

## <span data-ttu-id="c1b38-159"><a id="resource-tokens"></a>Pomocí minimální životnosti tokenu pro generovaných tokenů prostředků</span><span class="sxs-lookup"><span data-stu-id="c1b38-159"><a id="resource-tokens"></a>Use minimum token lifetimes for generated Resource tokens</span></span>

| <span data-ttu-id="c1b38-160">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-160">Title</span></span>                   | <span data-ttu-id="c1b38-161">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-161">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-162">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-162">**Component**</span></span>               | <span data-ttu-id="c1b38-163">Azure Documentdb</span><span class="sxs-lookup"><span data-stu-id="c1b38-163">Azure Document DB</span></span> | 
| <span data-ttu-id="c1b38-164">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-164">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-165">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-165">Build</span></span> |  
| <span data-ttu-id="c1b38-166">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-166">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-167">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-167">Generic</span></span> |
| <span data-ttu-id="c1b38-168">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-168">**Attributes**</span></span>              | <span data-ttu-id="c1b38-169">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-169">N/A</span></span>  |
| <span data-ttu-id="c1b38-170">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-170">**References**</span></span>              | <span data-ttu-id="c1b38-171">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-171">N/A</span></span>  |
| <span data-ttu-id="c1b38-172">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-172">**Steps**</span></span> | <span data-ttu-id="c1b38-173">Snižte časový interval tokenu prostředků na minimální požadovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c1b38-173">Reduce the timespan of resource token to a minimum value required.</span></span> <span data-ttu-id="c1b38-174">Prostředek tokeny mají platný časový interval výchozí 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="c1b38-174">Resource tokens have a default valid timespan of 1 hour.</span></span>|

## <span data-ttu-id="c1b38-175"><a id="wsfederation-logout"></a>Implementace správné odhlášení metodami WsFederation při použití služby AD FS</span><span class="sxs-lookup"><span data-stu-id="c1b38-175"><a id="wsfederation-logout"></a>Implement proper logout using WsFederation methods when using ADFS</span></span>

| <span data-ttu-id="c1b38-176">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-176">Title</span></span>                   | <span data-ttu-id="c1b38-177">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-177">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-178">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-178">**Component**</span></span>               | <span data-ttu-id="c1b38-179">ADFS</span><span class="sxs-lookup"><span data-stu-id="c1b38-179">ADFS</span></span> | 
| <span data-ttu-id="c1b38-180">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-180">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-181">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-181">Build</span></span> |  
| <span data-ttu-id="c1b38-182">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-182">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-183">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-183">Generic</span></span> |
| <span data-ttu-id="c1b38-184">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-184">**Attributes**</span></span>              | <span data-ttu-id="c1b38-185">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-185">N/A</span></span>  |
| <span data-ttu-id="c1b38-186">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-186">**References**</span></span>              | <span data-ttu-id="c1b38-187">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-187">N/A</span></span>  |
| <span data-ttu-id="c1b38-188">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-188">**Steps**</span></span> | <span data-ttu-id="c1b38-189">Pokud aplikace spoléhá na službu tokenů zabezpečení tokenem vydaným pomocí služby AD FS, obslužné rutiny události odhlášení by měly volat metodu WSFederationAuthenticationModule.FederatedSignOut() pro odhlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="c1b38-189">If the application relies on STS token issued by ADFS, the logout event handler should call WSFederationAuthenticationModule.FederatedSignOut() method to log out the user.</span></span> <span data-ttu-id="c1b38-190">Také musí být zničený aktuální relace a hodnota tokenu relace by měl resetování a zrušeny.</span><span class="sxs-lookup"><span data-stu-id="c1b38-190">Also the current session should be destroyed, and the session token value should be reset and nullified.</span></span>|

### <a name="example"></a><span data-ttu-id="c1b38-191">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-191">Example</span></span>
```C#
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes the user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at the specified security token service (STS) by using the WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of the current session and raises the appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at the specified security token service (STS) by using the WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <span data-ttu-id="c1b38-192"><a id="proper-logout"></a>Implementace správné odhlášení při použití serveru identit</span><span class="sxs-lookup"><span data-stu-id="c1b38-192"><a id="proper-logout"></a>Implement proper logout when using Identity Server</span></span>

| <span data-ttu-id="c1b38-193">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-193">Title</span></span>                   | <span data-ttu-id="c1b38-194">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-194">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-195">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-195">**Component**</span></span>               | <span data-ttu-id="c1b38-196">Serveru identit</span><span class="sxs-lookup"><span data-stu-id="c1b38-196">Identity Server</span></span> | 
| <span data-ttu-id="c1b38-197">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-197">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-198">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-198">Build</span></span> |  
| <span data-ttu-id="c1b38-199">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-199">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-200">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-200">Generic</span></span> |
| <span data-ttu-id="c1b38-201">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-201">**Attributes**</span></span>              | <span data-ttu-id="c1b38-202">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-202">N/A</span></span>  |
| <span data-ttu-id="c1b38-203">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-203">**References**</span></span>              | [<span data-ttu-id="c1b38-204">Federované IdentityServer3 odhlášení</span><span class="sxs-lookup"><span data-stu-id="c1b38-204">IdentityServer3-Federated sign out</span></span>](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| <span data-ttu-id="c1b38-205">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-205">**Steps**</span></span> | <span data-ttu-id="c1b38-206">IdentityServer podporuje možnost vytvořit federaci s poskytovateli externí identity.</span><span class="sxs-lookup"><span data-stu-id="c1b38-206">IdentityServer supports the ability to federate with external identity providers.</span></span> <span data-ttu-id="c1b38-207">Když uživatel odhlásí od zprostředkovatele identity nadřazeného, v závislosti na použitém protokolu, může být obdržet oznámení, když se uživatel odhlásí. To umožňuje IdentityServer k upozornění klientů, jeho, takže se můžete také odhlášení uživatele. Podívejte se do dokumentace v části odkazy podrobnosti implementace.</span><span class="sxs-lookup"><span data-stu-id="c1b38-207">When a user signs out of an upstream identity provider, depending upon the protocol used, it might be possible to receive a notification when the user signs out. It allows IdentityServer to notify its clients so they can also sign the user out. Check the documentation in the references section for the implementation details.</span></span>|

## <span data-ttu-id="c1b38-208"><a id="https-secure-cookies"></a>Aplikace dostupné přes protokol HTTPS, musíte použít bezpečné soubory cookie</span><span class="sxs-lookup"><span data-stu-id="c1b38-208"><a id="https-secure-cookies"></a>Applications available over HTTPS must use secure cookies</span></span>

| <span data-ttu-id="c1b38-209">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-209">Title</span></span>                   | <span data-ttu-id="c1b38-210">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-210">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-211">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-211">**Component**</span></span>               | <span data-ttu-id="c1b38-212">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-212">Web Application</span></span> | 
| <span data-ttu-id="c1b38-213">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-213">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-214">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-214">Build</span></span> |  
| <span data-ttu-id="c1b38-215">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-215">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-216">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-216">Generic</span></span> |
| <span data-ttu-id="c1b38-217">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-217">**Attributes**</span></span>              | <span data-ttu-id="c1b38-218">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="c1b38-218">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="c1b38-219">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-219">**References**</span></span>              | <span data-ttu-id="c1b38-220">[Element (schéma nastavení ASP.NET) httpCookies](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure vlastnost](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span><span class="sxs-lookup"><span data-stu-id="c1b38-220">[httpCookies Element (ASP.NET Settings Schema)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure Property](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span></span> |
| <span data-ttu-id="c1b38-221">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-221">**Steps**</span></span> | <span data-ttu-id="c1b38-222">Soubory cookie jsou obvykle přístupné pouze k doméně, pro kterou byly obor.</span><span class="sxs-lookup"><span data-stu-id="c1b38-222">Cookies are normally only accessible to the domain for which they were scoped.</span></span> <span data-ttu-id="c1b38-223">Bohužel definici "domény" nezahrnuje protokol, soubory cookie, které jsou vytvořené pomocí protokolu HTTPS jsou přístupné přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1b38-223">Unfortunately, the definition of "domain" does not include the protocol so cookies that are created over HTTPS are accessible over HTTP.</span></span> <span data-ttu-id="c1b38-224">Atribut "zabezpečení" označuje do prohlížeče, soubor cookie měl pouze být k dispozici prostřednictvím protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c1b38-224">The "secure" attribute indicates to the browser that the cookie should only be made available over HTTPS.</span></span> <span data-ttu-id="c1b38-225">Zkontrolujte, že všechny soubory cookie nastavená přes HTTPS pomocí **zabezpečené** atribut.</span><span class="sxs-lookup"><span data-stu-id="c1b38-225">Ensure that all cookies set over HTTPS use the **secure** attribute.</span></span> <span data-ttu-id="c1b38-226">Požadavek na lze vynutit v souboru web.config nastavením requireSSL atributu na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="c1b38-226">The requirement can be enforced in the web.config file by setting the requireSSL attribute to true.</span></span> <span data-ttu-id="c1b38-227">Je žádoucí, protože se vynutit **zabezpečené** atribut pro všechny soubory cookie aktuálních a budoucích bez nutnosti žádné změny další kód.</span><span class="sxs-lookup"><span data-stu-id="c1b38-227">It is the preferred approach because it will enforce the **secure** attribute for all current and future cookies without the need to make any additional code changes.</span></span>|

### <a name="example"></a><span data-ttu-id="c1b38-228">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-228">Example</span></span>
```C#
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
<span data-ttu-id="c1b38-229">Nastavení se nevynucuje i v případě, že HTTP se používá pro přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c1b38-229">The setting is enforced even if HTTP is used to access the application.</span></span> <span data-ttu-id="c1b38-230">Pokud HTTP se používá pro přístup k aplikaci, dělí nastavení aplikace, protože soubory cookie se nastavují s atribut zabezpečení a prohlížeče nebude přidán do aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1b38-230">If HTTP is used to access the application, the setting breaks the application because the cookies are set with the secure attribute and the browser will not send them back to the application.</span></span>

| <span data-ttu-id="c1b38-231">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-231">Title</span></span>                   | <span data-ttu-id="c1b38-232">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-232">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-233">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-233">**Component**</span></span>               | <span data-ttu-id="c1b38-234">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-234">Web Application</span></span> | 
| <span data-ttu-id="c1b38-235">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-235">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-236">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-236">Build</span></span> |  
| <span data-ttu-id="c1b38-237">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-237">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-238">Webové formuláře, MVC5</span><span class="sxs-lookup"><span data-stu-id="c1b38-238">Web Forms, MVC5</span></span> |
| <span data-ttu-id="c1b38-239">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-239">**Attributes**</span></span>              | <span data-ttu-id="c1b38-240">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="c1b38-240">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="c1b38-241">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-241">**References**</span></span>              | <span data-ttu-id="c1b38-242">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-242">N/A</span></span>  |
| <span data-ttu-id="c1b38-243">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-243">**Steps**</span></span> | <span data-ttu-id="c1b38-244">Pokud webová aplikace je předávající strana a rozšíření IdP je ADFS server, nastavením requireSSL na hodnotu True v je možné nakonfigurovat zabezpečené atribut tokenu FedAuth `system.identityModel.services` části souboru Web.config:</span><span class="sxs-lookup"><span data-stu-id="c1b38-244">When the web application is the Relying Party, and the IdP is ADFS server, the FedAuth token's secure attribute can be configured by setting requireSSL to True in `system.identityModel.services` section of web.config:</span></span>|

### <a name="example"></a><span data-ttu-id="c1b38-245">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-245">Example</span></span>
```C#
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <span data-ttu-id="c1b38-246"><a id="cookie-definition"></a>Všechny aplikace http, na základě by měl určovat pouze pro definici souboru cookie http</span><span class="sxs-lookup"><span data-stu-id="c1b38-246"><a id="cookie-definition"></a>All http based application should specify http only for cookie definition</span></span>

| <span data-ttu-id="c1b38-247">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-247">Title</span></span>                   | <span data-ttu-id="c1b38-248">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-248">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-249">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-249">**Component**</span></span>               | <span data-ttu-id="c1b38-250">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-250">Web Application</span></span> | 
| <span data-ttu-id="c1b38-251">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-251">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-252">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-252">Build</span></span> |  
| <span data-ttu-id="c1b38-253">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-253">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-254">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-254">Generic</span></span> |
| <span data-ttu-id="c1b38-255">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-255">**Attributes**</span></span>              | <span data-ttu-id="c1b38-256">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-256">N/A</span></span>  |
| <span data-ttu-id="c1b38-257">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-257">**References**</span></span>              | [<span data-ttu-id="c1b38-258">Zabezpečený soubor Cookie atribut</span><span class="sxs-lookup"><span data-stu-id="c1b38-258">Secure Cookie Attribute</span></span>](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| <span data-ttu-id="c1b38-259">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-259">**Steps**</span></span> | <span data-ttu-id="c1b38-260">Chcete-li zmírnit riziko zpřístupnění informací s útoku skriptování (XSS), nový atribut - httpOnly - zavedl se soubory cookie a podporuje všechny hlavní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="c1b38-260">To mitigate the risk of information disclosure with a cross-site scripting (XSS) attack, a new attribute - httpOnly - was introduced to cookies and is supported by all major browsers.</span></span> <span data-ttu-id="c1b38-261">Atribut určuje, že soubor cookie není přístupná prostřednictvím skriptu.</span><span class="sxs-lookup"><span data-stu-id="c1b38-261">The attribute specifies that a cookie is not accessible through script.</span></span> <span data-ttu-id="c1b38-262">Webovou aplikaci pomocí souborů cookie HttpOnly snižuje možnost, můžete důvěrné informace obsažené v souboru cookie odcizení pomocí skriptu a posílá webu útočníka.</span><span class="sxs-lookup"><span data-stu-id="c1b38-262">By using HttpOnly cookies, a web application reduces the possibility that sensitive information contained in the cookie can be stolen via script and sent to an attacker's website.</span></span> |

### <a name="example"></a><span data-ttu-id="c1b38-263">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-263">Example</span></span>
<span data-ttu-id="c1b38-264">Všechny aplikace založené na protokolu HTTP, které používají soubory cookie by měl v definici souboru cookie zadejte HttpOnly implementací následující konfigurace v souboru web.config:</span><span class="sxs-lookup"><span data-stu-id="c1b38-264">All HTTP-based applications that use cookies should specify HttpOnly in the cookie definition, by implementing following configuration in web.config:</span></span>
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| <span data-ttu-id="c1b38-265">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-265">Title</span></span>                   | <span data-ttu-id="c1b38-266">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-266">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-267">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-267">**Component**</span></span>               | <span data-ttu-id="c1b38-268">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-268">Web Application</span></span> | 
| <span data-ttu-id="c1b38-269">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-269">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-270">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-270">Build</span></span> |  
| <span data-ttu-id="c1b38-271">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-271">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-272">Webové formuláře</span><span class="sxs-lookup"><span data-stu-id="c1b38-272">Web Forms</span></span> |
| <span data-ttu-id="c1b38-273">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-273">**Attributes**</span></span>              | <span data-ttu-id="c1b38-274">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-274">N/A</span></span>  |
| <span data-ttu-id="c1b38-275">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-275">**References**</span></span>              | [<span data-ttu-id="c1b38-276">Vlastnost FormsAuthentication.RequireSSL</span><span class="sxs-lookup"><span data-stu-id="c1b38-276">FormsAuthentication.RequireSSL Property</span></span>](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| <span data-ttu-id="c1b38-277">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-277">**Steps**</span></span> | <span data-ttu-id="c1b38-278">Hodnota vlastnosti RequireSSL je nastavena v konfiguračním souboru aplikace ASP.NET pomocí atributu requireSSL prvku konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c1b38-278">The RequireSSL property value is set in the configuration file for an ASP.NET application by using the requireSSL attribute of the configuration element.</span></span> <span data-ttu-id="c1b38-279">Zadaný v souboru Web.config vaší aplikace zda vrátit souboru cookie ověřování založené na formulářích na server nastavením atributu requireSSL je požadován protokol SSL (Secure Sockets Layer).</span><span class="sxs-lookup"><span data-stu-id="c1b38-279">You can specify in the Web.config file for your ASP.NET application whether SSL (Secure Sockets Layer) is required to return the forms-authentication cookie to the server by setting the requireSSL attribute.</span></span>|

### <a name="example"></a><span data-ttu-id="c1b38-280">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-280">Example</span></span> 
<span data-ttu-id="c1b38-281">Následující příklad kódu nastaví atribut requireSSL v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="c1b38-281">The following code example sets the requireSSL attribute in the Web.config file.</span></span>
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| <span data-ttu-id="c1b38-282">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-282">Title</span></span>                   | <span data-ttu-id="c1b38-283">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-283">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-284">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-284">**Component**</span></span>               | <span data-ttu-id="c1b38-285">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-285">Web Application</span></span> | 
| <span data-ttu-id="c1b38-286">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-286">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-287">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-287">Build</span></span> |  
| <span data-ttu-id="c1b38-288">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-288">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-289">MVC5</span><span class="sxs-lookup"><span data-stu-id="c1b38-289">MVC5</span></span> |
| <span data-ttu-id="c1b38-290">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-290">**Attributes**</span></span>              | <span data-ttu-id="c1b38-291">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="c1b38-291">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="c1b38-292">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-292">**References**</span></span>              | [<span data-ttu-id="c1b38-293">Windows Identity Foundation (WIF) konfigurace – část II</span><span class="sxs-lookup"><span data-stu-id="c1b38-293">Windows Identity Foundation (WIF) Configuration – Part II</span></span>](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| <span data-ttu-id="c1b38-294">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-294">**Steps**</span></span> | <span data-ttu-id="c1b38-295">Pokud chcete nastavit atribut httpOnly pro soubory cookie FedAuth, hideFromCsript atribut je třeba nastavit na hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="c1b38-295">To set httpOnly attribute for FedAuth cookies, hideFromCsript attribute value should be set to True.</span></span> |

### <a name="example"></a><span data-ttu-id="c1b38-296">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-296">Example</span></span>
<span data-ttu-id="c1b38-297">Následující konfigurace zobrazuje správné konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="c1b38-297">Following configuration shows the correct configuration:</span></span>
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

## <span data-ttu-id="c1b38-298"><a id="csrf-asp"></a>Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na webových stránkách ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c1b38-298"><a id="csrf-asp"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>

| <span data-ttu-id="c1b38-299">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-299">Title</span></span>                   | <span data-ttu-id="c1b38-300">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-300">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-301">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-301">**Component**</span></span>               | <span data-ttu-id="c1b38-302">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-302">Web Application</span></span> | 
| <span data-ttu-id="c1b38-303">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-303">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-304">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-304">Build</span></span> |  
| <span data-ttu-id="c1b38-305">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-305">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-306">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-306">Generic</span></span> |
| <span data-ttu-id="c1b38-307">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-307">**Attributes**</span></span>              | <span data-ttu-id="c1b38-308">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-308">N/A</span></span>  |
| <span data-ttu-id="c1b38-309">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-309">**References**</span></span>              | <span data-ttu-id="c1b38-310">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-310">N/A</span></span>  |
| <span data-ttu-id="c1b38-311">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-311">**Steps**</span></span> | <span data-ttu-id="c1b38-312">Padělání požadavku posílaného mezi weby (proti útokům CSRF nebo XSRF) je typ útoku, ve kterém může útočník provádět akce v kontextu zabezpečení navázanou relaci jiného uživatele na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="c1b38-312">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in the security context of a different user's established session on a web site.</span></span> <span data-ttu-id="c1b38-313">Cílem je upravit nebo odstranit obsah, pokud cílové webu závisí výhradně na soubory cookie relace k ověření přijatý požadavek.</span><span class="sxs-lookup"><span data-stu-id="c1b38-313">The goal is to modify or delete content, if the targeted web site relies exclusively on session cookies to authenticate received request.</span></span> <span data-ttu-id="c1b38-314">Útočník by mohl zneužít tuto chybu zabezpečení tím, že získáme prohlížeče jiný uživatel se načíst adresu URL pomocí příkazu z citlivé lokality, na kterém je již přihlášen.</span><span class="sxs-lookup"><span data-stu-id="c1b38-314">An attacker could exploit this vulnerability by getting a different user's browser to load a URL with a command from a vulnerable site on which the user is already logged in.</span></span> <span data-ttu-id="c1b38-315">Existuje mnoho způsobů pro útočníka na to, jako třeba hostování na jiný web, který načítá prostředku ze serveru citlivé nebo získávání uživatele klikněte na odkaz.</span><span class="sxs-lookup"><span data-stu-id="c1b38-315">There are many ways for an attacker to do that, such as by hosting a different web site that loads a resource from the vulnerable server, or getting the user to click a link.</span></span> <span data-ttu-id="c1b38-316">Pokud server odešle klientovi další token, vyžaduje, aby klient mají být zahrnuty všechny budoucí požadavky tento token a ověřuje, že všechny budoucí požadavky patří token, který se týká aktuální relaci, například pomocí technologie ASP.NET se dá zabránit útokům AntiForgeryToken nebo stavu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c1b38-316">The attack can be prevented if the server sends an additional token to the client, requires the client to include that token in all future requests, and verifies that all future requests include a token that pertains to the current session, such as by using the ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="c1b38-317">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-317">Title</span></span>                   | <span data-ttu-id="c1b38-318">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-318">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-319">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-319">**Component**</span></span>               | <span data-ttu-id="c1b38-320">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-320">Web Application</span></span> | 
| <span data-ttu-id="c1b38-321">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-321">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-322">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-322">Build</span></span> |  
| <span data-ttu-id="c1b38-323">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-323">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-324">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="c1b38-324">MVC5, MVC6</span></span> |
| <span data-ttu-id="c1b38-325">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-325">**Attributes**</span></span>              | <span data-ttu-id="c1b38-326">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-326">N/A</span></span>  |
| <span data-ttu-id="c1b38-327">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-327">**References**</span></span>              | [<span data-ttu-id="c1b38-328">Prevence XSRF/proti útokům CSRF v architektuře ASP.NET MVC a webových stránek</span><span class="sxs-lookup"><span data-stu-id="c1b38-328">XSRF/CSRF Prevention in ASP.NET MVC and Web Pages</span></span>](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| <span data-ttu-id="c1b38-329">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-329">**Steps**</span></span> | <span data-ttu-id="c1b38-330">Anti-proti útokům CSRF a formulářů rozhraní ASP.NET MVC – použít `AntiForgeryToken` pomocnou metodu pro zobrazení; put `Html.AntiForgeryToken()` do formátu, například</span><span class="sxs-lookup"><span data-stu-id="c1b38-330">Anti-CSRF and ASP.NET MVC forms - Use the `AntiForgeryToken` helper method on Views; put an `Html.AntiForgeryToken()` into the form, for example,</span></span>|

### <a name="example"></a><span data-ttu-id="c1b38-331">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-331">Example</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a><span data-ttu-id="c1b38-332">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-332">Example</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="c1b38-333">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-333">Example</span></span>
<span data-ttu-id="c1b38-334">Ve stejnou dobu Html.AntiForgeryToken() dává návštěvníka souboru cookie s názvem __RequestVerificationToken se stejnou hodnotou jako náhodné hodnoty hidden uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="c1b38-334">At the same time, Html.AntiForgeryToken() gives the visitor a cookie called __RequestVerificationToken, with the same value as the random hidden value shown above.</span></span> <span data-ttu-id="c1b38-335">V dalším kroku k ověření příchozí post formuláře, přidejte filtr [ValidateAntiForgeryToken] do cílové metody akce.</span><span class="sxs-lookup"><span data-stu-id="c1b38-335">Next, to validate an incoming form post, add the [ValidateAntiForgeryToken] filter to the target action method.</span></span> <span data-ttu-id="c1b38-336">Například:</span><span class="sxs-lookup"><span data-stu-id="c1b38-336">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="c1b38-337">Filtr autorizace, který kontroluje, zda:</span><span class="sxs-lookup"><span data-stu-id="c1b38-337">Authorization filter that checks that:</span></span>
* <span data-ttu-id="c1b38-338">Příchozí požadavek má souboru cookie s názvem __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="c1b38-338">The incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="c1b38-339">Příchozí požadavek má `Request.Form` položka názvem __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="c1b38-339">The incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="c1b38-340">Tyto soubory cookie a `Request.Form` za předpokladu, že všechny shody hodnoty je dobře, žádost prochází jako normální.</span><span class="sxs-lookup"><span data-stu-id="c1b38-340">These cookie and `Request.Form` values match Assuming all is well, the request goes through as normal.</span></span> <span data-ttu-id="c1b38-341">Ale pokud ne, pak chybu autorizace se zprávou "požadovaný token proti padělání nebyl zadán nebo byl neplatný".</span><span class="sxs-lookup"><span data-stu-id="c1b38-341">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span> 

### <a name="example"></a><span data-ttu-id="c1b38-342">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-342">Example</span></span>
<span data-ttu-id="c1b38-343">Ochrana proti útokům CSRF a AJAX: tokenu formuláře může být problém pro požadavky AJAX, protože požadavek AJAX může odesílat data JSON, ne data formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="c1b38-343">Anti-CSRF and AJAX: The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="c1b38-344">Jedno řešení je odeslat tokenů v vlastní hlavičku HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1b38-344">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="c1b38-345">Následující kód používá syntaxi Razor ke generování tokenů a potom přidá tokenů na požadavek AJAX.</span><span class="sxs-lookup"><span data-stu-id="c1b38-345">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> 
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

### <a name="example"></a><span data-ttu-id="c1b38-346">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-346">Example</span></span>
<span data-ttu-id="c1b38-347">Při zpracovávání požadavku extrahuje tokeny z hlaviček požadavku.</span><span class="sxs-lookup"><span data-stu-id="c1b38-347">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="c1b38-348">Potom zavolejte metodu AntiForgery.Validate k ověřování tokenů.</span><span class="sxs-lookup"><span data-stu-id="c1b38-348">Then call the AntiForgery.Validate method to validate the tokens.</span></span> <span data-ttu-id="c1b38-349">Metodu Validate vyvolá výjimku, pokud tokeny nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="c1b38-349">The Validate method throws an exception if the tokens are not valid.</span></span>
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

| <span data-ttu-id="c1b38-350">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-350">Title</span></span>                   | <span data-ttu-id="c1b38-351">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-351">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-352">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-352">**Component**</span></span>               | <span data-ttu-id="c1b38-353">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-353">Web Application</span></span> | 
| <span data-ttu-id="c1b38-354">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-354">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-355">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-355">Build</span></span> |  
| <span data-ttu-id="c1b38-356">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-356">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-357">Webové formuláře</span><span class="sxs-lookup"><span data-stu-id="c1b38-357">Web Forms</span></span> |
| <span data-ttu-id="c1b38-358">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-358">**Attributes**</span></span>              | <span data-ttu-id="c1b38-359">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-359">N/A</span></span>  |
| <span data-ttu-id="c1b38-360">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-360">**References**</span></span>              | [<span data-ttu-id="c1b38-361">Využití výhod funkcí ASP.NET integrované do jim útoky webové</span><span class="sxs-lookup"><span data-stu-id="c1b38-361">Take Advantage of ASP.NET Built-in Features to Fend Off Web Attacks</span></span>](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| <span data-ttu-id="c1b38-362">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-362">**Steps**</span></span> | <span data-ttu-id="c1b38-363">Útoky proti útokům CSRF v aplikacích webový formulář na základě omezeny nastavení ViewStateUserKey náhodný řetězec, který se liší pro jednotlivé uživatele – ID uživatele, nebo ještě lepší ID relace.</span><span class="sxs-lookup"><span data-stu-id="c1b38-363">CSRF attacks in WebForm based applications can be mitigated by setting ViewStateUserKey to a random string that varies for each user - user ID or, better yet, session ID.</span></span> <span data-ttu-id="c1b38-364">Pro počtu technická a sociálních důvodů ID relace je mnohem lepší umístit, protože relace na ID nepředvídatelným, časový limit a liší se na jednotlivé uživatele.</span><span class="sxs-lookup"><span data-stu-id="c1b38-364">For a number of technical and social reasons, session ID is a much better fit because a session ID is unpredictable, times out, and varies on a per-user basis.</span></span>|

### <a name="example"></a><span data-ttu-id="c1b38-365">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-365">Example</span></span>
<span data-ttu-id="c1b38-366">Tady je kód, který je potřeba mít v všechny stránky:</span><span class="sxs-lookup"><span data-stu-id="c1b38-366">Here's the code you need to have in all of your pages:</span></span>
```C#
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <span data-ttu-id="c1b38-367"><a id="inactivity-lifetime"></a>Nastavení relace dobu jeho existence nečinnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-367"><a id="inactivity-lifetime"></a>Set up session for inactivity lifetime</span></span>

| <span data-ttu-id="c1b38-368">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-368">Title</span></span>                   | <span data-ttu-id="c1b38-369">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-369">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-370">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-370">**Component**</span></span>               | <span data-ttu-id="c1b38-371">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-371">Web Application</span></span> | 
| <span data-ttu-id="c1b38-372">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-372">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-373">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-373">Build</span></span> |  
| <span data-ttu-id="c1b38-374">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-374">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-375">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-375">Generic</span></span> |
| <span data-ttu-id="c1b38-376">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-376">**Attributes**</span></span>              | <span data-ttu-id="c1b38-377">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-377">N/A</span></span>  |
| <span data-ttu-id="c1b38-378">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-378">**References**</span></span>              | <span data-ttu-id="c1b38-379">[Vlastnost HttpSessionState.Timeout](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="c1b38-379">[HttpSessionState.Timeout Property](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span></span> |
| <span data-ttu-id="c1b38-380">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-380">**Steps**</span></span> | <span data-ttu-id="c1b38-381">Časový limit relace představuje událostí, ke kterým dochází, když uživatel neprovede žádnou akci na webovém serveru během intervalu (definovanou webový server).</span><span class="sxs-lookup"><span data-stu-id="c1b38-381">Session timeout represents the event occurring when a user does not perform any action on a web site during a interval (defined by web server).</span></span> <span data-ttu-id="c1b38-382">Událost na straně serveru, změňte stav relace uživatele na "neplatný. (například" nepoužívá už") a na server WWW zrušení jeho (odstranění všech dat obsažených do ní).</span><span class="sxs-lookup"><span data-stu-id="c1b38-382">The event, on server side, change the status of the user session to 'invalid' (for example  "not used anymore") and instruct the web server to destroy it (deleting all data contained into it).</span></span> <span data-ttu-id="c1b38-383">Následující příklad kódu nastaví atribut časový limit relace na 15 minut v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="c1b38-383">The following code example sets the timeout session attribute to 15 minutes in the Web.config file.</span></span>|

### <a name="example"></a><span data-ttu-id="c1b38-384">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-384">Example</span></span>
<span data-ttu-id="c1b38-385">"" Kód XML <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration></span><span class="sxs-lookup"><span data-stu-id="c1b38-385">``\`XML code <configuration> <system.web> <sessionState mode="InProc" cookieless="true" timeout="15" /> </system.web> </configuration></span></span>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| <span data-ttu-id="c1b38-386">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-386">Title</span></span>                   | <span data-ttu-id="c1b38-387">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-387">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-388">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-388">**Component**</span></span>               | <span data-ttu-id="c1b38-389">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-389">Web Application</span></span> | 
| <span data-ttu-id="c1b38-390">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-390">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-391">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-391">Build</span></span> |  
| <span data-ttu-id="c1b38-392">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-392">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-393">Webové formuláře</span><span class="sxs-lookup"><span data-stu-id="c1b38-393">Web Forms</span></span> |
| <span data-ttu-id="c1b38-394">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-394">**Attributes**</span></span>              | <span data-ttu-id="c1b38-395">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-395">N/A</span></span>  |
| <span data-ttu-id="c1b38-396">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-396">**References**</span></span>              | <span data-ttu-id="c1b38-397">[forms Element pro ověřování (schéma nastavení ASP.NET)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="c1b38-397">[forms Element for authentication (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span></span> |
| <span data-ttu-id="c1b38-398">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-398">**Steps**</span></span> | <span data-ttu-id="c1b38-399">Nastavit časový limit souboru cookie lístek pro ověřování pomocí formulářů na 15 minut</span><span class="sxs-lookup"><span data-stu-id="c1b38-399">Set the Forms Authentication Ticket cookie timeout to 15 minutes</span></span>|

### <a name="example"></a><span data-ttu-id="c1b38-400">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-400">Example</span></span>
<span data-ttu-id="c1b38-401">"" Kód XML<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span><span class="sxs-lookup"><span data-stu-id="c1b38-401">``\`XML code <forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span></span>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When the web application is Relying Party and ADFS is the STS, the lifetime of the authentication cookies - FedAuth tokens - can be set by the following configuration in web.config:|

### Example
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use the code below to enable encryption-decryption of claims received from ADFS. Thumbprint value varies based on the certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a><span data-ttu-id="c1b38-402">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-402">Example</span></span>
<span data-ttu-id="c1b38-403">Také služby AD FS vydán SAML deklarace identity, že doba platnosti tokenu musí být nastavena na 15 minut, a to spuštěním následujícího příkazu powershellu na serveru služby AD FS:</span><span class="sxs-lookup"><span data-stu-id="c1b38-403">Also the ADFS issued SAML claim token's lifetime should be set to 15 minutes, by executing the following powershell command on the ADFS server:</span></span>
```C#
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <span data-ttu-id="c1b38-404"><a id="proper-app-logout"></a>Implementace správné odhlášení z aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-404"><a id="proper-app-logout"></a>Implement proper logout from the application</span></span>

| <span data-ttu-id="c1b38-405">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-405">Title</span></span>                   | <span data-ttu-id="c1b38-406">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-406">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-407">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-407">**Component**</span></span>               | <span data-ttu-id="c1b38-408">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c1b38-408">Web Application</span></span> | 
| <span data-ttu-id="c1b38-409">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-409">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-410">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-410">Build</span></span> |  
| <span data-ttu-id="c1b38-411">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-411">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-412">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-412">Generic</span></span> |
| <span data-ttu-id="c1b38-413">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-413">**Attributes**</span></span>              | <span data-ttu-id="c1b38-414">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-414">N/A</span></span>  |
| <span data-ttu-id="c1b38-415">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-415">**References**</span></span>              | <span data-ttu-id="c1b38-416">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-416">N/A</span></span>  |
| <span data-ttu-id="c1b38-417">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-417">**Steps**</span></span> | <span data-ttu-id="c1b38-418">Proveďte správné odhlásit z aplikace, pokud uživatele stisknutí odhlášení tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c1b38-418">Perform proper Sign Out from the application, when user presses log out button.</span></span> <span data-ttu-id="c1b38-419">Po odhlášení aplikace by měl destroy uživatelské relace a také resetování a nezruší se tím hodnota souboru cookie relace, spolu s obnovením a anulovány hodnota souboru cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="c1b38-419">Upon logout, application should destroy user's session, and also reset and nullify session cookie value, along with resetting and nullifying authentication cookie value.</span></span> <span data-ttu-id="c1b38-420">Navíc pokud více relací, jsou svázané s identitou jednoho uživatele, se musí být souhrnně ukončena na straně serveru při vypršení časového limitu nebo odhlášení.</span><span class="sxs-lookup"><span data-stu-id="c1b38-420">Also, when multiple sessions are tied to a single user identity, they must be collectively terminated on the server side at timeout or logout.</span></span> <span data-ttu-id="c1b38-421">Nakonec zkontrolujte, že odhlášení funkce je k dispozici na každé stránce.</span><span class="sxs-lookup"><span data-stu-id="c1b38-421">Lastly, ensure that Logout functionality is available on every page.</span></span> |

## <span data-ttu-id="c1b38-422"><a id="csrf-api"></a>Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c1b38-422"><a id="csrf-api"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>

| <span data-ttu-id="c1b38-423">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-423">Title</span></span>                   | <span data-ttu-id="c1b38-424">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-424">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-425">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-425">**Component**</span></span>               | <span data-ttu-id="c1b38-426">Web API</span><span class="sxs-lookup"><span data-stu-id="c1b38-426">Web API</span></span> | 
| <span data-ttu-id="c1b38-427">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-427">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-428">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-428">Build</span></span> |  
| <span data-ttu-id="c1b38-429">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-429">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-430">Obecné</span><span class="sxs-lookup"><span data-stu-id="c1b38-430">Generic</span></span> |
| <span data-ttu-id="c1b38-431">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-431">**Attributes**</span></span>              | <span data-ttu-id="c1b38-432">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-432">N/A</span></span>  |
| <span data-ttu-id="c1b38-433">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-433">**References**</span></span>              | <span data-ttu-id="c1b38-434">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-434">N/A</span></span>  |
| <span data-ttu-id="c1b38-435">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-435">**Steps**</span></span> | <span data-ttu-id="c1b38-436">Padělání požadavku posílaného mezi weby (proti útokům CSRF nebo XSRF) je typ útoku, ve kterém může útočník provádět akce v kontextu zabezpečení navázanou relaci jiného uživatele na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="c1b38-436">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in the security context of a different user's established session on a web site.</span></span> <span data-ttu-id="c1b38-437">Cílem je upravit nebo odstranit obsah, pokud cílové webu závisí výhradně na soubory cookie relace k ověření přijatý požadavek.</span><span class="sxs-lookup"><span data-stu-id="c1b38-437">The goal is to modify or delete content, if the targeted web site relies exclusively on session cookies to authenticate received request.</span></span> <span data-ttu-id="c1b38-438">Útočník by mohl zneužít tuto chybu zabezpečení tím, že získáme prohlížeče jiný uživatel se načíst adresu URL pomocí příkazu z citlivé lokality, na kterém je již přihlášen.</span><span class="sxs-lookup"><span data-stu-id="c1b38-438">An attacker could exploit this vulnerability by getting a different user's browser to load a URL with a command from a vulnerable site on which the user is already logged in.</span></span> <span data-ttu-id="c1b38-439">Existuje mnoho způsobů pro útočníka na to, jako třeba hostování na jiný web, který načítá prostředku ze serveru citlivé nebo získávání uživatele klikněte na odkaz.</span><span class="sxs-lookup"><span data-stu-id="c1b38-439">There are many ways for an attacker to do that, such as by hosting a different web site that loads a resource from the vulnerable server, or getting the user to click a link.</span></span> <span data-ttu-id="c1b38-440">Pokud server odešle klientovi další token, vyžaduje, aby klient mají být zahrnuty všechny budoucí požadavky tento token a ověřuje, že všechny budoucí požadavky patří token, který se týká aktuální relaci, například pomocí technologie ASP.NET se dá zabránit útokům AntiForgeryToken nebo stavu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c1b38-440">The attack can be prevented if the server sends an additional token to the client, requires the client to include that token in all future requests, and verifies that all future requests include a token that pertains to the current session, such as by using the ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="c1b38-441">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-441">Title</span></span>                   | <span data-ttu-id="c1b38-442">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-442">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-443">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-443">**Component**</span></span>               | <span data-ttu-id="c1b38-444">Web API</span><span class="sxs-lookup"><span data-stu-id="c1b38-444">Web API</span></span> | 
| <span data-ttu-id="c1b38-445">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-445">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-446">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-446">Build</span></span> |  
| <span data-ttu-id="c1b38-447">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-447">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-448">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="c1b38-448">MVC5, MVC6</span></span> |
| <span data-ttu-id="c1b38-449">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-449">**Attributes**</span></span>              | <span data-ttu-id="c1b38-450">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c1b38-450">N/A</span></span>  |
| <span data-ttu-id="c1b38-451">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-451">**References**</span></span>              | [<span data-ttu-id="c1b38-452">Prevence útoků (proti útokům CSRF) padělání požadavku posílaného mezi weby v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c1b38-452">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| <span data-ttu-id="c1b38-453">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-453">**Steps**</span></span> | <span data-ttu-id="c1b38-454">Ochrana proti útokům CSRF a AJAX: tokenu formuláře může být problém pro požadavky AJAX, protože požadavek AJAX může odesílat data JSON, ne data formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="c1b38-454">Anti-CSRF and AJAX: The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="c1b38-455">Jedno řešení je odeslat tokenů v vlastní hlavičku HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1b38-455">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="c1b38-456">Následující kód používá syntaxi Razor ke generování tokenů a potom přidá tokenů na požadavek AJAX.</span><span class="sxs-lookup"><span data-stu-id="c1b38-456">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> |

### <a name="example"></a><span data-ttu-id="c1b38-457">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-457">Example</span></span>
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

### <a name="example"></a><span data-ttu-id="c1b38-458">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-458">Example</span></span>
<span data-ttu-id="c1b38-459">Při zpracovávání požadavku extrahuje tokeny z hlaviček požadavku.</span><span class="sxs-lookup"><span data-stu-id="c1b38-459">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="c1b38-460">Potom zavolejte metodu AntiForgery.Validate k ověřování tokenů.</span><span class="sxs-lookup"><span data-stu-id="c1b38-460">Then call the AntiForgery.Validate method to validate the tokens.</span></span> <span data-ttu-id="c1b38-461">Metodu Validate vyvolá výjimku, pokud tokeny nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="c1b38-461">The Validate method throws an exception if the tokens are not valid.</span></span>
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

### <a name="example"></a><span data-ttu-id="c1b38-462">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-462">Example</span></span>
<span data-ttu-id="c1b38-463">Anti-proti útokům CSRF a formulářů rozhraní ASP.NET MVC – využít pomocnou metodu AntiForgeryToken na zobrazení; Uveďte Html.AntiForgeryToken() do formuláře, například</span><span class="sxs-lookup"><span data-stu-id="c1b38-463">Anti-CSRF and ASP.NET MVC forms - Use the AntiForgeryToken helper method on Views; put an Html.AntiForgeryToken() into the form, for example,</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a><span data-ttu-id="c1b38-464">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-464">Example</span></span>
<span data-ttu-id="c1b38-465">V předchozím příkladu bude výstup přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="c1b38-465">The example above will output something like the following:</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="c1b38-466">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-466">Example</span></span>
<span data-ttu-id="c1b38-467">Ve stejnou dobu Html.AntiForgeryToken() dává návštěvníka souboru cookie s názvem __RequestVerificationToken se stejnou hodnotou jako náhodné hodnoty hidden uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="c1b38-467">At the same time, Html.AntiForgeryToken() gives the visitor a cookie called __RequestVerificationToken, with the same value as the random hidden value shown above.</span></span> <span data-ttu-id="c1b38-468">V dalším kroku k ověření příchozí post formuláře, přidejte filtr [ValidateAntiForgeryToken] do cílové metody akce.</span><span class="sxs-lookup"><span data-stu-id="c1b38-468">Next, to validate an incoming form post, add the [ValidateAntiForgeryToken] filter to the target action method.</span></span> <span data-ttu-id="c1b38-469">Například:</span><span class="sxs-lookup"><span data-stu-id="c1b38-469">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="c1b38-470">Filtr autorizace, který kontroluje, zda:</span><span class="sxs-lookup"><span data-stu-id="c1b38-470">Authorization filter that checks that:</span></span>
* <span data-ttu-id="c1b38-471">Příchozí požadavek má souboru cookie s názvem __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="c1b38-471">The incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="c1b38-472">Příchozí požadavek má `Request.Form` položka názvem __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="c1b38-472">The incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="c1b38-473">Tyto soubory cookie a `Request.Form` za předpokladu, že všechny shody hodnoty je dobře, žádost prochází jako normální.</span><span class="sxs-lookup"><span data-stu-id="c1b38-473">These cookie and `Request.Form` values match Assuming all is well, the request goes through as normal.</span></span> <span data-ttu-id="c1b38-474">Ale pokud ne, pak chybu autorizace se zprávou "požadovaný token proti padělání nebyl zadán nebo byl neplatný".</span><span class="sxs-lookup"><span data-stu-id="c1b38-474">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span>

| <span data-ttu-id="c1b38-475">Název</span><span class="sxs-lookup"><span data-stu-id="c1b38-475">Title</span></span>                   | <span data-ttu-id="c1b38-476">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c1b38-476">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="c1b38-477">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="c1b38-477">**Component**</span></span>               | <span data-ttu-id="c1b38-478">Web API</span><span class="sxs-lookup"><span data-stu-id="c1b38-478">Web API</span></span> | 
| <span data-ttu-id="c1b38-479">**SDL fáze**</span><span class="sxs-lookup"><span data-stu-id="c1b38-479">**SDL Phase**</span></span>               | <span data-ttu-id="c1b38-480">Sestavení</span><span class="sxs-lookup"><span data-stu-id="c1b38-480">Build</span></span> |  
| <span data-ttu-id="c1b38-481">**Použít technologie**</span><span class="sxs-lookup"><span data-stu-id="c1b38-481">**Applicable Technologies**</span></span> | <span data-ttu-id="c1b38-482">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="c1b38-482">MVC5, MVC6</span></span> |
| <span data-ttu-id="c1b38-483">**Atributy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-483">**Attributes**</span></span>              | <span data-ttu-id="c1b38-484">Poskytovatel Identity identity zprostředkovatel – AD FS, – Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1b38-484">Identity Provider - ADFS, Identity Provider - Azure AD</span></span> |
| <span data-ttu-id="c1b38-485">**Odkazy**</span><span class="sxs-lookup"><span data-stu-id="c1b38-485">**References**</span></span>              | [<span data-ttu-id="c1b38-486">Zabezpečení webového rozhraní API s jednotlivých účtů a místní přihlášení v rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="c1b38-486">Secure a Web API with Individual Accounts and Local Login in ASP.NET Web API 2.2</span></span>](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| <span data-ttu-id="c1b38-487">**Kroky**</span><span class="sxs-lookup"><span data-stu-id="c1b38-487">**Steps**</span></span> | <span data-ttu-id="c1b38-488">Pokud webového rozhraní API je zabezpečené pomocí OAuth 2.0, pak se očekává nosný token v hlavičce autorizace požadavku a udělí přístup k požadavek pouze v případě, že token je platný.</span><span class="sxs-lookup"><span data-stu-id="c1b38-488">If the Web API is secured using OAuth 2.0, then it expects a bearer token in Authorization request header and grants access to the request only if the token is valid.</span></span> <span data-ttu-id="c1b38-489">Na rozdíl od ověřování na základě souborů cookie prohlížeče nepřipojujte nosné tokeny požadavků.</span><span class="sxs-lookup"><span data-stu-id="c1b38-489">Unlike cookie based authentication, browsers do not attach the bearer tokens to requests.</span></span> <span data-ttu-id="c1b38-490">Klienta, který je potřeba explicitně připojte nosný token v hlavičce žádosti.</span><span class="sxs-lookup"><span data-stu-id="c1b38-490">The requesting client needs to explicitly attach the bearer token in the request header.</span></span> <span data-ttu-id="c1b38-491">Pro rozhraní ASP.NET Web API chráněné pomocí OAuth 2.0, proto jsou nosné tokeny za obrana proti útokům proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="c1b38-491">Therefore, for ASP.NET Web APIs protected using OAuth 2.0, bearer tokens are considered as a defense against CSRF attacks.</span></span> <span data-ttu-id="c1b38-492">Upozorňujeme, že pokud část aplikace MVC používá ověřování pomocí formulářů (tj, soubory cookie používá), tokeny proti zfalšování musí být použity ve webové aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="c1b38-492">Please note that if the MVC portion of the application uses forms authentication (i.e., uses cookies), anti-forgery tokens have to be used by the MVC web app.</span></span> |

### <a name="example"></a><span data-ttu-id="c1b38-493">Příklad</span><span class="sxs-lookup"><span data-stu-id="c1b38-493">Example</span></span>
<span data-ttu-id="c1b38-494">Rozhraní Web API musí být informováni spolehnout jenom na nosné tokeny a ne v souborech cookie.</span><span class="sxs-lookup"><span data-stu-id="c1b38-494">The Web API has to be informed to rely ONLY on bearer tokens and not on cookies.</span></span> <span data-ttu-id="c1b38-495">Je možné ji provést následující konfigurace v `WebApiConfig.Register` metoda: '''C ostré kód konfigurace. SuppressDefaultHostAuthentication(); konfigurace. Filters.Add (nové HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span><span class="sxs-lookup"><span data-stu-id="c1b38-495">It can be done by the following configuration in `WebApiConfig.Register` method: ``\`C-Sharp code config.SuppressDefaultHostAuthentication(); config.Filters.Add(new HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span></span>
```
The SuppressDefaultHostAuthentication method tells Web API to ignore any authentication that happens before the request reaches the Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API to authenticate only using bearer tokens.